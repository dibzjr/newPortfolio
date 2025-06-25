<script>
import feather from 'feather-icons';
import ProjectHeader from '../components/projects/ProjectHeader.vue';
import ProjectGallery from '../components/projects/ProjectGallery.vue';
import ProjectInfo from '../components/projects/ProjectInfo.vue';
import ProjectRelatedProjects from '../components/projects/ProjectRelatedProjects.vue';

export default {
	name: 'Projects',
	components: {
		ProjectHeader,
		ProjectGallery,
		ProjectInfo,
		ProjectRelatedProjects,
	},
	data: () => {
		return {
			singleProjectHeader: {
				singleProjectTitle: 'Merchant Portal',
				singleProjectDate: 'Feb 26, 2025',
				singleProjectTag: 'UI / Frontend',
			},
			projectImages: [
				{
					id: 1,
					title: 'Kabul Project Management UI',
					img: require('@/assets/images/ui-project-1.jpg'),
				},
				{
					id: 2,
					title: 'Kabul Project Management UI',
					img: require('@/assets/images/web-project-2.jpg'),
				},
				{
					id: 3,
					title: 'Kabul Project Management UI',
					img: require('@/assets/images/mobile-project-2.jpg'),
				},
			],
			projectInfo: {
				clientHeading: 'About Client',
				companyInfos: [
					{
						id: 1,
						title: 'Name',
						details: 'DPO Pay',
					},
					{
						id: 2,
						title: 'Services',
						details: 'Payment Services',
					},
					{
						id: 3,
						title: 'Website',
						details: 'https://dpogroup.com/',
					},
					{
						id: 4,
						title: 'Project Link',
						details: 'https://f2f-portal.dpodev.biz',
					},
				],
				objectivesHeading: 'Objective',
				objectivesDetails:
					'Enable Single Sign On for nominated DPO Group merchants',
				technologies: [
					{
						title: 'Tools & Technologies',
						techs: [
							'HTML',
							'CSS',
							'TypeScript',
							'JavaScript',
							'Vue.js',
							'TailwindCSS',
							'PHP',
							'MSSQL',
							'MySql',
							'Golang',
							'Lambda',
							'High Charts'
						],
					},
				],
				projectDetailsHeading: 'Description',
				projectDetails: [
					{
						id: 1,
						details:
							'The project entails creating a visually appealing Merchant Portal with functionalities such as transaction creation and direct charging using cards and mobile, report fetching, data visualization using high charts js showcasing data such as sales, settlements, POS transactions e.t.c. ',
					},
					// {
					// 	id: 2,
					// 	details:
					// 		'With this relationship, the parent company is able to perform specific actions on behalf of the child account such as transaction creation, refunds, view details of child accounts such as balances e.t.c',
					// },
					// {
					// 	id: 3,
					// 	details:
					// 		'Lorem ipsum dolor, sit amet consectetur adipisicing elit. Nihil vel illum asperiores dignissimos cumque quibusdam et fugiat voluptatem nobis suscipit explicabo, eaque consequatur nesciunt, fugit eligendi corporis laudantium adipisci soluta?',
					// },
					// {
					// 	id: 4,
					// 	details:
					// 		'Lorem ipsum dolor, sit amet consectetur adipisicing elit. Nihil vel illum asperiores dignissimos cumque quibusdam et fugiat voluptatem nobis suscipit explicabo, eaque consequatur nesciunt, fugit eligendi corporis laudantium adipisci soluta? Lorem ipsum, dolor sit amet consectetur adipisicing elit. Incidunt totam dolorum, ducimus obcaecati, voluptas facilis molestias nobis ut quam natus similique inventore excepturi optio ipsa deleniti fugit illo. Unde, amet! Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ipsum illo necessitatibus perspiciatis! Aperiam perferendis labore temporibus, eos culpa corporis recusandae quas, fuga voluptatibus nesciunt odit libero tenetur neque consequatur ea.',
					// },
				],
				// socialSharingsHeading: 'Share This',
				// socialSharings: [
				// 	{
				// 		id: 1,
				// 		name: 'Twitter',
				// 		icon: 'twitter',
				// 		url: 'https://twitter.com/realstoman',
				// 	},
				// 	{
				// 		id: 2,
				// 		name: 'Instagram',
				// 		icon: 'instagram',
				// 		url: 'https://instagram.com/realstoman',
				// 	},
				// 	{
				// 		id: 3,
				// 		name: 'Facebook',
				// 		icon: 'facebook',
				// 		url: 'https://facebook.com/',
				// 	},
				// 	{
				// 		id: 4,
				// 		name: 'LinkedIn',
				// 		icon: 'linkedin',
				// 		url: 'https://linkedin.com/',
				// 	},
				// 	{
				// 		id: 5,
				// 		name: 'Youtube',
				// 		icon: 'youtube',
				// 		url: 'https://www.youtube.com/c/StomanStudio',
				// 	},
				// ],
			},
			relatedProject: {
				relatedProjectsHeading: 'Related Projects',
				relatedProjects: [
					{
						id: 1,
						title: 'Group Accounts',
						img: require('@/assets/images/web-project-2.jpg'),
						route: '/projects/single-project'
					},
					// {
					// 	id: 2,
					// 	title: 'Mobile UI',
					// 	img: require(' @/assets/images/mobile-project-1.jpg'),
					// },
					// {
					// 	id: 3,
					// 	title: 'UI Design',
					// 	img: require('@/assets/images/ui-project-2.jpg'),
					// },
					// {
					// 	id: 4,
					// 	title: 'Kabul Mobile App UI',
					// 	img: require('@/assets/images/mobile-project-2.jpg'),
					// },
				],
			},
		};
	},
	mounted() {
		feather.replace();
	},
	updated() {
		feather.replace();
	},
	methods: {},
};
</script>

<template>
	<div class="container mx-auto mt-10 sm:mt-20">
		<!-- Project header -->
		<ProjectHeader :singleProjectHeader="singleProjectHeader" />

		<!-- Project gallery -->
		<ProjectGallery :projectImages="projectImages" />

		<!-- Project information -->
		<ProjectInfo :projectInfo="projectInfo" />

		<!-- Project related projects -->
		<ProjectRelatedProjects :relatedProject="relatedProject" />
	</div>
</template>

<style scoped></style>
